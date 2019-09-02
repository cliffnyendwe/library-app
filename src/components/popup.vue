<template>
    <v-dialog max-width="600px" max-height="1200px" v-model="dialog">
       <v-chip flat slot="activator" class="teal darken-3 white--text">Add new Book</v-chip>
        <v-card>
            <v-card-author>
                <h2>Add a new Book</h2>
            </v-card-author>
            <v-card-text>
                <v-form  ref="form">
                    <v-text-field label="Book author" :rules="inputrules" v-model="author" prepend-icon="folder"></v-text-field>
                     <v-textarea v-model="description" :rules="inputrules"             label="Book Details" prepend-icon="edit"></v-textarea>
                        <v-menu>
                            <v-text-field   :value="formattedDate" slot="activator" label="Due date" prepend-icon="date_range"></v-text-field>
                                <v-date-picker v-model="due"></v-date-picker>

                        </v-menu>
                        <v-spacer></v-spacer>
                        <v-btn flat @click="submit" :loading="loading" class="teal mx-5 mt-3">Add Book</v-btn>
                </v-form>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>

import format from 'date-fns/format'

export default {
    data(){
        return{
            author:'',
            description:'',
            borrow-status: '',
            availability: '',
            due: null,
            loading: false,
            dialog:false
    }
},
  methods: {
        submit(){
            if(this.$refs.form.validate()){
                this.loading=true;

                const book ={
                    author:this.author,
                    description:this.description,
                    borrow-status:this.borrow-status,
                    availability:this.availability,
                    due:format(this.due,'Do MMMM YYYY'),
                    book: 'User',
                    status: 'completed'


                }

                db.collection('books').add(book).then(()=>{

                    this.loading=false;
                    this.dialog=false;
                    this.$emit('bookadded')
                })
            }

        },
    },
    computed:{
        formattedDate(){

            return this.due ? format(this.due, 'Do MMM YYYY'):''
        }
    }
}
</script>
