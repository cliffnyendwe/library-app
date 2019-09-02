<template>
    <div class="dashboard">
        <h1 class="subheading grey--text"><v-icon>dashboard</v-icon>Dashboard</h1>
            <v-container class="my-5">
                <v-layout row class="mb-3">
                    <v-tooltip top>
                        <v-btn small flat color="grey" slot="activator">
                            <v-icon small left>folder</v-icon>
                            <span class="caption text-lowercase" @click="sortBy('author')">By book Name</span>
                        </v-btn>
                              <span>Sort books by book name</span>
                    </v-tooltip>

                     <v-tooltip top>
                        <v-btn small flat color="grey" slot="activator">
                            <v-icon small left>book</v-icon>
                            <span class="caption text-lowercase" @click="sortBy('book')">By book Name</span>
                        </v-btn>
                        <span>Sort books by book</span>
                      </v-tooltip>
                </v-layout>
                <v-card flat class="pa-5" v-for="book in books" :key="book.authore">
                    <!-- Using typescript 6 so as to give styling according to a property in the array i.e the status property  -->
                    <v-layout row wrap :class="`pa-3 book ${book.status}`">
                        <v-flex xs12 md6>
                           <div class="caption grey--text">book authore</div>
                           <div>{{book.authore}}</div>
                        </v-flex>
                        <v-flex xs6 sm4 md2>
                           <div class="caption grey--text">book</div>
                            <div>{{book.book}}</div>
                        </v-flex>
                         <v-flex xs6 sm4 md2>
                           <div class="caption grey--text">Due By</div>
                            <div>{{book.due}}</div>
                        </v-flex>
                        <v-flex xs2 sm4 md2>
                            <div class="right">
                                <v-chip small :class="`${book.status} white--text my-2 caption`">{{ book.status }}</v-chip>
                            </div>
                        </v-flex>
                    </v-layout>

                </v-card>
            </v-container>
    </div>
</template>

<script>
import db from '@/fb'
export default {
    data(){
    return {
      books: []
    }
  },
  methods:{
      sortBy(prop){
        //  Sort takes a callback funtion with two parameters compares the two values
          this.books.sort((a,b)=>a[prop]< b[prop] ? -1:1)
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
<style>

.book.complete{
  border-left: 4px solid #3CD1C2;
}
.book.ongoing{
  border-left: 4px solid orange
}
.book.overdue{
  border-left: 4px solid tomato;
}
.v-chip.complete{
  background: #3cd1c2;
}
.v-chip.ongoing{
  background: #ffaa2c;
}
.v-chip.overdue{
  background: #f83e70;
}

</style>
