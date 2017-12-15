<template>
  <div class="hello">
<!--  Input user  -->
    <div class="columns is-mobile is-centered">
      <div class="column is-half is-narrow">
        <p class="bd-notification is-info">

          <div class="level-item">
            <div class="field has-addons">
              <p class="control">
                <input class="input" type="text"  v-model="data.Name"  placeholder="Name">
              </p>
              <p class="control">
                <input class="input" type="text"  v-model="data.Surname" placeholder="Surname">
              </p>
              <p class="control">
                <input class="input" type="number"  v-model="data.Phone" placeholder="Phone">
              </p>
              <p class="control">
                <button class="button is-primary is-outlined"  @click="add()" >
                Insert
                </button>
              </p>

            </div>
          </div>

    </p>
  </div>
</div>
<!--  จบ Input user  -->

<!--  ตาราง user  -->
<div class="columns is-mobile is-centered">
  <div class="column is-half is-narrow">
    <p class="bd-notification is-info">

      <div  class="table1" >
          <table class="table">
                <thead>
                  <tr>
                    <th scope="col">#</th>
                    <th scope="col">Name</th>
                    <th scope="col">Surname</th>
                    <th scope="col">Phone</th>
                    <th scope="col">update</th>
                    <th scope="col">Delete Account</th>
                  </tr>
                </thead>

                  <tbody v-if="checkEdit !== key" v-for = " (User, key, count) in showUser">
                    <tr>
                      <td>  {{count+1}}  </td>
                      <td> {{User.Name}} </td>
                      <td> {{User.Surname}}  </td>
                      <td> {{User.Phone}}  </td>
                       <td> <button type="button" class="button is-primary is-outlined" name="buttonAdd" @click="swap(key)" >update</button> </td>
                      <td>
                        <a class="button is-danger is-outlined"><span  @click="remove(key)" >Delete</span><span class="icon is-small">  <i class="fa fa-times"></i></span></a>
                      </td>
                    </tr>
                  </tbody>

                  <tbody class="content" v-else>
                    <tr>
                      <td>  {{count+1}}  </td>
                      <td> <input class="input" type="text"  v-model="data.Name"  > </td>
                      <td> <input class="input" type="text"  v-model="data.Surname" >  </td>
                      <td> <input class="input" type="number"  v-model="data.Phone" >  </td>
                      <td> <button type="button" class="button is-info is-outlined" name="buttonAdd" @click="update(key,data.Name,data.Surname,data.Phone)" >Approved</button> </td>
                    </tr>
                  </tbody>
          </table>
      </div>

    </p>
  </div>
</div>
<!-- จบ ตาราง user  -->

  </div>
</template>

<script>
// Initialize Firebaseง
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyC9YLdXomMdwGEHhpCGe8eGF49R4-HKFik',
  authDomain: 'finaltestvue.firebaseapp.com',
  databaseURL: 'https://finaltestvue.firebaseio.com',
  projectId: 'finaltestvue',
  storageBucket: 'finaltestvue.appspot.com',
  messagingSenderId: '556891722962'
}
firebase.initializeApp(config)
export default {
  name: 'HelloWorld',
  data () {
    return {
      data: {
        Name: '',
        Surname: '',
        Phone: ''
      },
      showUser: '',
      checkEdit: ''
    }
  },
  created: function () {
    this.pullData()
  },
  methods: {
    pullData: function () {  /* แสดงชืตาราง Admin */
      let that = this
      firebase.database().ref('/User/').once('value').then(function (snapshot) {
        that.showUser = snapshot.val()
      })
    },
    add: function () {
      firebase.database().ref('User').push(this.data)
      alert('Add Admin Complete')
      this.pullData()
      this.data = ''
    },
    update: function (key, Name, Surname, Phone) {
      console.log(key)
      alert('Add Credit Complete')
      firebase.database().ref('/User/').child(key).update({
        Name: Name,
        Phone: Phone,
        Surname: Surname
      })
      this.checkEdit = ''
      this.pullData()
    },
    swap: function (key) {
      this.checkEdit = key
    },
    remove: function (key) {
      alert('Delete Admin Complete')
      firebase.database().ref('User').child(key).remove()
      this.pullData()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
