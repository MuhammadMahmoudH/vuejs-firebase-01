<template>
    <div id="app" class="container">
        <div class="page-header">
            <h1>Vue.js 2 & Firebase <small>Sample Application by CodingTheSmartWay.com</small></h1>
        </div>
        <div class="panel panel-default">
            <div class="panel-heading">
                <h3 class="panel-title">Add New Books</h3>
            </div>
            <div class="panel-body">
                <form id="form" class="form-inline" v-on:submit.prevent="addBook">
                    <div class="form-group">
                        <label for="bookTitle">Title:</label>
                        <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
                    </div>
                    <div class="form-group">
                        <label for="bookAuthor">Author:</label>
                        <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
                    </div>
                    <div class="form-group">
                        <label for="bookUrl">Url:</label>
                        <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
                    </div>
                    <input type="submit" class="btn btn-primary" value="Add Book">
                </form>
            </div>
        </div>
        <div class="panel panel-default">
            <div class="panel-heading">
                <h3 class="panel-title">Book List</h3>
            </div>
            <div class="panel-body">
                <table class="table table-striped">
                    <thead>
                    <tr>
                        <th>Title</th>
                        <th>Author</th>
                        <th></th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="book in books">
                        <td><a v-bind:href="book.url">{{book.title}}</a></td>
                        <td>{{book.author}}</td>
                        <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span></td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>

    import Firebase from 'firebase'

    import toastr from 'toastr'

    let config = {
        apiKey: "AIzaSyBuB7rTFxdk5l0cL8w8-hdkBLXVst1xqEI",
        authDomain: "vue-firebase-01-ea639.firebaseapp.com",
        databaseURL: "https://vue-firebase-01-ea639.firebaseio.com",
        projectId: "vue-firebase-01-ea639",
        storageBucket: "vue-firebase-01-ea639.appspot.com",
        messagingSenderId: "46524891689"
    };

    let app = Firebase.initializeApp(config)
    let db = app.database()

    let booksRef = db.ref('books')


    export default {
        name: 'app',

        firebase: {
            books: booksRef
        },

        data () {
            return {
                newBook: {
                    title: '',
                    author: '',
                    url: 'http://'
                }
            }
        },

        methods: {
            addBook(){
                booksRef.push(this.newBook);
                this.newBook.title = '';
                this.newBook.author = '';
                this.newBook.url = 'http://';
            },
            removeBook(book){
                booksRef.child(book['.key']).remove()
                toastr.success('تم الحذف بنجاح', 'سوق.كوم',{showMethod : 'slideDown'},{hideMethod : 'slideUp'}, {timeOut: 5000})

            }
        },



    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        margin-top: 20px;
    }
</style>