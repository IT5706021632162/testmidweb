<template>
  <div class="hello">
    <div class="columns">
            <div class="column"></div>
                <div class="column is-two-thirds">
                  <div class="columns">
                    <div class="column">
                      <div class="field">
                        <div class="control">
                          <input class="input is-info" type="number"  v-model="code" placeholder="รหัสนักศึกษา">
                        </div>
                      </div>
                    </div>

                    <div class="column">
                      <div class="field">
                        <div class="control">
                          <input class="input is-info" type="text" v-model="name" placeholder="ชื่อ">
                        </div>
                      </div>
                    </div>

                    <div class="column">
                      <div class="field">
                          <div class="control">
                            <input class="input is-info" type="text" v-model="surname" placeholder="นามสกุล">
                          </div>
                        </div>
                    </div>

                    <div class="column">
                      <a class="button is-info is-outlined" @click="insert()">Add</a>
                    </div>
                </div>
              </div>
            <div class="column"></div>
          </div>
          <br>

          <div class="columns">
            <div class="column"></div>
              <div class="column is-two-thirds">
                <div  class="tablestudent" >
                  <table class="table" >
                    <thead>
                      <tr>
                        <th scope="col">#</th>
                        <th scope="col">Code</th>
                        <th scope="col">Name</th>
                        <th scope="col">Surname</th>
                      </tr>
                    </thead>
                    <tbody   v-if="checkEdit !== key" v-for = "(show, key, count) in showstudent"  :key ="show.num">
                      <tr>
                        <td>  {{count+1}}  </td>
                        <td>  {{show.code}}  </td>
                        <td>  {{show.name}} </td>
                        <td>  {{show.surname}} </td>
                        <td>      <a class="button is-info is-outlined"  @click="swap(key)">Update</a> </td>
                        <td>      <a class="button is-info is-outlined" @click="Delete(key)">Delete</a> </td>
                      </tr>
                    </tbody>
                    <tbody v-else >
                      <tr >
                         <td>  {{count+1}}  </td>
                          <td><input class="input is-info" type="number"  v-model="show.code" placeholder="รหัสนักศึกษา"> </td>
                        <td>  <input class="input is-info" type="text" v-model="show.name" placeholder="ชื่อ"> </td>
                        <td>  <input class="input is-info" type="text" v-model="show.surname" placeholder="นามสกุล"> </td>
                        <td> <button type="button" class="button button is-success " name="buttonAdd"  @click="Update(key,show.code,show.name,show.surname)" >Save</button> </td>
                        <td> <button type="button" class="button is-danger" name="buttonAdd" @click="cancel()" >Cancel</button> </td>

                      </tr>
                    </tbody>
                  </table>
              </div>
            </div>
          </div>
  </div>
</template>

<script>
import firebase from 'firebase'
// Initialize Firebase
var config = {
  apiKey: 'AIzaSyCjl88WLHmFMuCgyfsTdHiibOLVJV9FIrs',
  authDomain: 'testmud-36590.firebaseapp.com',
  databaseURL: 'https://testmud-36590.firebaseio.com',
  projectId: 'testmud-36590',
  storageBucket: 'testmud-36590.appspot.com',
  messagingSenderId: '1097521811911'
}
firebase.initializeApp(config)
export default {
  name: 'HelloWorld',
  data () {
    return {
      showstudent: '',
      cout: 0,
      checkEdit: '',
      data: {
        code: '',
        name: '',
        surname: ''
      }
    }
  },
  created: function () {
    this.pullData()
  },
  methods: {
    pullData: function () {
      let that = this
      firebase.database().ref('/student/').once('value').then(function (snapshot) {
        that.showstudent = snapshot.val()
      })
    },
    insert () {
      this.data.code = this.code
      this.data.name = this.name
      this.data.surname = this.surname
      firebase.database().ref('student').push(this.data)
      this.pullData()
    },
    Update (key, code, name, sur) {
      firebase.database().ref('/student/').child(key).update({
        code: code,
        name: name,
        surname: sur
      })
      this.pullData()
      this.checkEdit = ''
    },
    Delete (key) {
      firebase.database().ref('student').child(key).remove()
      this.pullData()
    },
    swap: function (key) {
      this.checkEdit = key
    },
    cancel () {
      this.checkEdit = ''
    }
  }
}
</script>
